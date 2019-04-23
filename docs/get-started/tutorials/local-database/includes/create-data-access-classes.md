---
ms.openlocfilehash: 83e28796a2c387927dddd708da3ee6623f800a35
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61193253"
---
In dieser Übung fügen Sie dem Projekt **LocalDatabaseTutorial** Datenzugriffsklassen hinzu, mit denen Personendaten in der Datenbank gespeichert werden.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Fügen Sie im **Projektmappen-Explorer** dem Projekt **LocalDatabaseTutorial** eine neue Klasse mit dem Namen `Person` hinzu. Entfernen Sie anschließend in **Person.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    In diesem Codeausschnitt wird eine `Person`-Klasse definiert, die Daten zu allen Personen in der Anwendung speichert. Die `ID`-Eigenschaft wird mit den Attributen `PrimaryKey` und `AutoIncrement` markiert, um sicherzustellen, dass jede `Person`-Instanz in der Datenbank über eine eindeutige ID verfügt, die von SQLite.NET vergeben wird.

1. Fügen Sie im **Projektmappen-Explorer** dem Projekt **LocalDatabaseTutorial** eine neue Klasse mit dem Namen `Database` hinzu. Entfernen Sie anschließend in **Database.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    Diese Klasse enthält Code, mit dem die Datenbank erstellt wird und Daten aus ihr gelesen bzw. in sie geschrieben werden. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `Database`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad wird in der nächsten Übung von der `App`-Klasse bereitgestellt.

1. Erweitern Sie **App.xaml** im **Projektmappen-Explorer** im Projekt **LocalDatabaseTutorial**, und doppelklicken Sie dann auf die Datei **App.xaml.cs**, um sie zu öffnen. Entfernen Sie anschließend in **App.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    In diesem Codeausschnitt wird eine `Database`-Eigenschaft definiert, mit der eine neue `Database`-Instanz als Singleton erstellt wird. Als Argumente für den Konstruktor der `Database`-Klasse werden ein lokaler Dateipfad und ein Dateiname als Speicherort der Datenbank übergeben.

    > [!IMPORTANT Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Fügen Sie im **Lösungspad** dem Projekt **LocalDatabaseTutorial** eine neue Klasse mit dem Namen `Person` hinzu. Entfernen Sie anschließend in **Person.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    In diesem Codeausschnitt wird eine `Person`-Klasse definiert, die Daten zu allen Personen in der Anwendung speichert. Die `ID`-Eigenschaft wird mit den Attributen `PrimaryKey` und `AutoIncrement` markiert, um sicherzustellen, dass jede `Person`-Instanz in der Datenbank über eine eindeutige ID verfügt, die von SQLite.NET vergeben wird.

1. Fügen Sie im **Lösungspad** dem Projekt **LocalDatabaseTutorial** eine neue Klasse mit dem Namen `Database` hinzu. Entfernen Sie anschließend in **Database.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    Diese Klasse enthält Code, mit dem die Datenbank erstellt wird und Daten aus ihr gelesen bzw. in sie geschrieben werden. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `Database`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad wird in der nächsten Übung von der `App`-Klasse bereitgestellt.

1. Erweitern Sie **App.xaml** im **Lösungspad** im Projekt **LocalDatabaseTutorial**, und doppelklicken Sie dann auf die Datei **App.xaml.cs**, um sie zu öffnen. Entfernen Sie anschließend in **App.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    In diesem Codeausschnitt wird eine `Database`-Eigenschaft definiert, mit der eine neue `Database`-Instanz als Singleton erstellt wird. Als Argumente für den Konstruktor der `Database`-Klasse werden ein lokaler Dateipfad und ein Dateiname als Speicherort der Datenbank übergeben.

    > [!IMPORTANT Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.
    
1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.
