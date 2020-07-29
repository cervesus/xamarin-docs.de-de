---
title: Speichern von Daten in einer lokalen SQLite.NET-Datenbank
description: In diesem Artikel erfahren Sie, wie Sie Daten in einer lokalen SQLite.NET-Datenbank speichern.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a28e391c6bacd460f095c94e30b2d9433a5191e4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931793"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>Speichern von Daten in einer lokalen SQLite.NET-Datenbank

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

In diesem Schnellstart lernen Sie, wie Sie:

- den NuGet-Paket-Manager verwenden, um ein NuGet-Paket zu einem Projekt hinzufügen.
- Daten lokal in einer SQLite.NET-Datenbank speichern.

In diesem Schnellstart erfahren Sie, wie Sie Daten in einer lokalen SQLite.NET-Datenbank speichern. Die fertige Anwendung wird unten gezeigt:

[![Notizenseite](database-images/screenshots1-sml.png)](database-images/screenshots1.png#lightbox "NotesPage")
[![Notizeneintragsseite](database-images/screenshots2-sml.png)](database-images/screenshots2.png#lightbox "NoteEntryPage")

## <a name="prerequisites"></a>Voraussetzungen

Sie sollten den [vorherigen Schnellstart](multi-page.md) erfolgreich abgeschlossen haben, bevor Sie mit diesem Schnellstart beginnen. Alternativ können Sie auch das [letzte Schnellstartbeispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/) herunterladen und als Startpunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio, und öffnen Sie die Projektmappe „Notizen“.

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Notizen** und anschließend auf **NuGet-Pakete verwalten...** :

    ![Hinzufügen von NuGet-Paketen](database-images/vs/add-nuget-packages.png)    

3. Klicken Sie im **NuGet-Paket-Manager** auf die Registerkarte **Durchsuchen**, suchen Sie nach dem NuGet-Paket **sqlite-net-pcl**, wählen Sie es aus, und klicken Sie auf die Schaltfläche **Installieren**, um es dem Projekt hinzuzufügen:

    ![Hinzufügen eines Pakets](database-images/vs/add-package.png)

    > [!NOTE]
    > Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:
    > - **Autor(en):** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Trotz des Paketnamens kann dieses NuGet-Paket in .NET Standard-Projekten verwendet werden.

    Mit diesem Paket können Datenbankvorgänge in der Anwendung verwendet werden.

4. Öffnen Sie im **Projektmappen-Explorer** im Projekt **Notizen** die Datei **Note.cs** im Ordner **Modelle**, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Diese Klasse definiert ein `Note`-Modell, das Daten über jede Notiz in der Anwendung speichert. Die `ID`-Eigenschaft wird mit den Attributen `PrimaryKey` und `AutoIncrement` markiert, um sicherzustellen, dass jede `Note`-Instanz in der SQLite.NET-Datenbank über eine eindeutige ID verfügt, die von SQLite.NET vergeben wird.

    Speichern Sie die Änderungen an **Note.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

5. Fügen Sie im **Projektmappen-Explorer** einen neuen Ordner namens **Daten** zum Projekt **Notizen** hinzu.

6. Fügen Sie im **Projektmappen-Explorer** im Projekt **Notizen** dem Ordner **Daten** eine neue Klasse mit dem Namen **NoteDatabase** hinzu.

7. Ersetzen Sie in **NoteDatabase.cs** den vorhandenen Code durch den folgenden Code:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    Diese Klasse enthält Code, mit dem die Datenbank erstellt wird und Daten aus ihr gelesen bzw. in sie geschrieben und aus ihr gelöscht werden. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `NoteDatabase`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad wird im nächsten Schritt von der `App`-Klasse bereitgestellt.

    Speichern Sie die Änderungen an **NoteDatabase.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

8. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notizen** auf **App.xaml.cs**, um die Datei zu öffnen. Ersetzen Sie dann den vorhandenen Code durch folgenden Code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
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

    Dieser Code definiert eine `Database`-Eigenschaft, die eine neue `NoteDatabase`-Instanz als Singleton erzeugt, wobei der Dateiname der Datenbank als Argument an den `NoteDatabase`-Konstruktor übergeben wird. Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

9. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notizen** auf **NotesPage.xaml.cs**, um diese Datei zu öffnen. Ersetzen Sie dann die `OnAppearing`-Methode durch folgenden Code:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Dieser Code befüllt die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse mit sämtlichen Notizen, die in der Datenbank gespeichert sind.

    Speichern Sie die Änderungen an **NotesPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

10. Doppelklicken Sie im **Projektmappen-Explorer** auf **NoteEntryPage.xaml.cs**, um diese Datei zu öffnen. Ersetzen Sie dann die `OnSaveButtonClicked`- und `OnDeleteButtonClicked`-Methoden durch folgenden Code:

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage` speichert eine `Note`-Instanz, die eine einzelne Notiz darstellt, in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite. Wenn der `OnSaveButtonClicked`-Ereignishandler ausgeführt wird, wird die `Note`-Instanz in der Datenbank gespeichert, und die Anwendung navigiert zur vorherigen Seite zurück. Wenn der `OnDeleteButtonClicked`-Ereignishandler ausgeführt wird, wird die `Note`-Instanz aus der Datenbank gelöscht, und die Anwendung navigiert zur vorherigen Seite zurück.

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

11. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie dieses aus. Weitere Informationen finden Sie unter [Erstellen des Schnellstarts](single-page.md#building-the-quickstart).

    Klicken Sie auf der **NotesPage**-Eigenschaft auf die Schaltfläche **+** , um zur Eigenschaft **NoteEntryPage** zu navigieren und eine Notiz einzugeben. Nach dem Speichern der Notiz navigiert die Anwendung zurück zur Eigenschaft **NotesPage**.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und öffnen Sie das Projekt „Notizen“.

2. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf das Projekt **Notizen** und anschließend auf **Add > Add NuGet Packages...** (Hinzufügen > NuGet-Pakete hinzufügen...):

    ![Hinzufügen von NuGet-Paketen](database-images/vsmac/add-nuget-packages.png)    

3. Suchen Sie im Fenster **Pakete hinzufügen** nach dem NuGet-Paket **sqlite-net-pcl**, wählen Sie es aus, und klicken Sie auf **Paket hinzufügen**, um es dem Projekt hinzuzufügen:

    ![Hinzufügen eines Pakets](database-images/vsmac/add-package.png)

    > [!NOTE]
    > Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:
    > - **Autor:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Trotz des Paketnamens kann dieses NuGet-Paket in .NET Standard-Projekten verwendet werden.

    Mit diesem Paket können Datenbankvorgänge in der Anwendung verwendet werden.

4. Öffnen Sie im **Lösungspad** im Projekt **Notizen** die Datei **Note.cs** im Ordner **Modelle**, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Diese Klasse definiert ein `Note`-Modell, das Daten über jede Notiz in der Anwendung speichert. Die `ID`-Eigenschaft wird mit den Attributen `PrimaryKey` und `AutoIncrement` markiert, um sicherzustellen, dass jede `Note`-Instanz in der SQLite.NET-Datenbank über eine eindeutige ID verfügt, die von SQLite.NET vergeben wird.

    Speichern Sie die Änderungen an **Note.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

5. Fügen Sie im **Lösungspad** einen neuen Ordner namens **Daten** zum Projekt **Notizen** hinzu.

6. Fügen Sie im **Lösungspad** im Projekt **Notizen** dem Ordner **Daten** eine neue Klasse mit dem Namen **NoteDatabase** hinzu.

7. Ersetzen Sie in **NoteDatabase.cs** den vorhandenen Code durch den folgenden Code:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    Diese Klasse enthält Code, mit dem die Datenbank erstellt wird und Daten aus ihr gelesen bzw. in sie geschrieben und aus ihr gelöscht werden. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `NoteDatabase`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad wird im nächsten Schritt von der `App`-Klasse bereitgestellt.

    Speichern Sie die Änderungen an **NoteDatabase.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

8. Doppelklicken Sie im **Lösungspad** im Projekt **Notizen** auf **App.xaml.cs**, um diese Datei zu öffnen. Ersetzen Sie dann den vorhandenen Code durch folgenden Code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
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

    Dieser Code definiert eine `Database`-Eigenschaft, die eine neue `NoteDatabase`-Instanz als Singleton erzeugt, wobei der Dateiname der Datenbank als Argument an den `NoteDatabase`-Konstruktor übergeben wird. Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

9. Doppelklicken Sie im **Lösungspad** im Projekt **Notizen** auf **NotesPage.xaml.cs**, um diese Datei zu öffnen. Ersetzen Sie dann die `OnAppearing`-Methode durch folgenden Code:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Dieser Code befüllt die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse mit sämtlichen Notizen, die in der Datenbank gespeichert sind.

    Speichern Sie die Änderungen an **NotesPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

10. Doppelklicken Sie im **Lösungspad** auf **NoteEntryPage.xaml.cs**, um die Datei zu öffnen. Ersetzen Sie dann die `OnSaveButtonClicked`- und `OnDeleteButtonClicked`-Methoden durch folgenden Code:

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage` speichert eine `Note`-Instanz, die eine einzelne Notiz darstellt, in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite. Wenn der `OnSaveButtonClicked`-Ereignishandler ausgeführt wird, wird die `Note`-Instanz in der Datenbank gespeichert, und die Anwendung navigiert zur vorherigen Seite zurück. Wenn der `OnDeleteButtonClicked`-Ereignishandler ausgeführt wird, wird die `Note`-Instanz aus der Datenbank gelöscht, und die Anwendung navigiert zur vorherigen Seite zurück.

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

11. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie dieses aus. Weitere Informationen finden Sie unter [Erstellen des Schnellstarts](single-page.md#building-the-quickstart).

    Klicken Sie auf der **NotesPage**-Eigenschaft auf die Schaltfläche **+** , um zur Eigenschaft **NoteEntryPage** zu navigieren und eine Notiz einzugeben. Nach dem Speichern der Notiz navigiert die Anwendung zurück zur Eigenschaft **NotesPage**.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie:

- den NuGet-Paket-Manager verwenden, um ein NuGet-Paket zu einem Projekt hinzufügen.
- Daten lokal in einer SQLite.NET-Datenbank speichern.

Fahren Sie mit dem nächsten Schnellstart fort, um die Anwendung mit XAML-Formatvorlagen zu gestalten.

> [!div class="nextstepaction"]
> [Nächste](styling.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md)
