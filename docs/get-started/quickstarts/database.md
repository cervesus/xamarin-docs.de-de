---
title: Speichern von Daten in einer lokalen SQLite.NET-Datenbank
description: In diesem Artikel wird erläutert, wie Sie Daten in einer lokalen sqlite.net-Datenbank speichern.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 2cd4726566e73aece5d0deef90ad1feedefaa2d8
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249683"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>Speichern von Daten in einer lokalen sqlite.net-Datenbank

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

In dieser Schnellstartanleitung erfahren Sie Folgendes:

- Verwenden Sie den nuget-Paket-Manager, um ein nuget-Paket zu einem Projekt hinzuzufügen.
- Lokale Speicherung von Daten in einer SQLite.net-Datenbank.

In der Schnellstartanleitung erfahren Sie, wie Sie Daten in einer lokalen sqlite.net-Datenbank speichern. Die fertige Anwendung wird unten gezeigt:

[![](database-images/screenshots1-sml.png "")](database-images/screenshots1.png#lightbox "Seite "Notizen"")
[Notiz![](database-images/screenshots2-sml.png "") der Seite "Notizen"](database-images/screenshots2.png#lightbox "Seite "Hinweis Eintrag"")

## <a name="prerequisites"></a>Erforderliche Komponenten

Sie sollten den [vorherigen Schnellstart](multi-page.md) erfolgreich abgeschlossen haben, bevor Sie diesen Schnellstart durchführen. Alternativ können Sie das [vorherige Schnellstart Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/) herunterladen und als Ausgangspunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio, und öffnen Sie die Lösung Notes.

2. Wählen Sie in **Projektmappen-Explorer** das Projekt **Notes** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **nuget-Pakete verwalten...** aus:

    ![](database-images/vs/add-nuget-packages.png "Hinzufügen von NuGet-Paketen")    

3. Klicken Sie im **NuGet-Paket-Manager** auf die Registerkarte **Durchsuchen**, suchen Sie nach dem NuGet-Paket **sqlite-net-pcl**, wählen Sie es aus, und klicken Sie auf die Schaltfläche **Installieren**, um es dem Projekt hinzuzufügen:

    ![](database-images/vs/add-package.png "Paket hinzufügen")

    > [!NOTE]
    > Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:
    > - **Autor(en):** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Trotz des Paketnamens kann dieses NuGet-Paket in .NET Standard-Projekten verwendet werden.

    Mit diesem Paket können Datenbankvorgänge in der Anwendung verwendet werden.

4. Öffnen Sie in **Projektmappen-Explorer** im Projekt " **Notizen** " **Note.cs** im Ordner " **Models** ", und ersetzen Sie den vorhandenen Code durch den folgenden Code:

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

    Diese Klasse definiert ein `Note` Modell, das Daten zu jeder Notiz in der Anwendung speichert. Die `ID` -Eigenschaft ist mit `PrimaryKey` dem `AutoIncrement` -Attribut und dem- `Note` Attribut gekennzeichnet, um sicherzustellen, dass jede Instanz in der SQLite.net-Datenbank eine eindeutige ID hat, die von SQLite.net

    Speichern Sie die Änderungen an **Note.cs** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

5. Fügen Sie in **Projektmappen-Explorer**dem **Notes** -Projekt einen neuen Ordner mit dem Namen " **Data** " hinzu.

6. Fügen Sie in **Projektmappen-Explorer**im Projekt **Notes** dem Ordner **Data** eine neue Klasse mit dem Namen **notedatabase** hinzu.

7. Ersetzen Sie in **NoteDatabase.cs**den vorhandenen Code durch den folgenden Code:

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

    Diese Klasse enthält Code zum Erstellen der Datenbank, zum Lesen von Daten, zum Schreiben von Daten in die Datenbank und zum Löschen von Daten aus der Datenbank. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `NoteDatabase`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad wird von der `App` -Klasse im nächsten Schritt bereitgestellt.

    Speichern Sie die Änderungen an **NoteDatabase.cs** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

8. Doppelklicken Sie in **Projektmappen-Explorer**im **Notes** -Projekt auf **app.XAML.cs** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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

    Dieser Code definiert eine `Database` Eigenschaft, die eine neue `NoteDatabase` Instanz als Singleton erstellt, wobei der Dateiname der Datenbank als Argument an den `NoteDatabase` Konstruktor übergeben wird. Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

9. Doppelklicken Sie in **Projektmappen-Explorer**im **Notes** -Projekt auf **NotesPage.XAML.cs** , um es zu öffnen. Ersetzen Sie anschließend `OnAppearing` die-Methode durch den folgenden Code:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Dieser Code füllt den [`ListView`](xref:Xamarin.Forms.ListView) mit allen in der Datenbank gespeicherten Notizen auf.

    Speichern Sie die Änderungen an **NotesPage.XAML.cs** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

10. Doppelklicken Sie in **Projektmappen-Explorer**auf **NoteEntryPage.XAML.cs** , um es zu öffnen. Ersetzen Sie anschließend `OnSaveButtonClicked` die `OnDeleteButtonClicked` -Methode und die-Methode durch den folgenden Code:

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

      Speichert eine `Note` -Instanz, die einen einzelnen Hinweis darstellt, in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite. `NoteEntryPage` Wenn der `OnSaveButtonClicked` Ereignishandler ausgeführt wird, wird `Note` die Instanz in der Datenbank gespeichert, und die Anwendung navigiert zurück zur vorherigen Seite. Wenn der `OnDeleteButtonClicked` Ereignishandler ausgeführt wird, wird `Note` die Instanz aus der Datenbank gelöscht, und die Anwendung navigiert zurück zur vorherigen Seite.

      Speichern Sie die Änderungen an **NoteEntryPage.XAML.cs** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

11. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter Erste Schritte mit [der Schnellstartanleitung](single-page.md#building-the-quickstart).

    Klicken Sie auf der **Seite NotesPage** auf die **+** Schaltfläche, um zur **noteentrypage** zu navigieren, und geben Sie einen Hinweis ein. Nach dem Speichern des Hinweises wird die Anwendung zurück zur **NotesPage**navigiert.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und öffnen Sie das Notes-Projekt.

2. Wählen Sie im **Lösungspad**das Projekt **Notes** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Hinzufügen > nuget-Pakete hinzufügen...** aus:

    ![](database-images/vsmac/add-nuget-packages.png "Hinzufügen von NuGet-Paketen")    

3. Suchen Sie im Fenster **Pakete hinzufügen** nach dem NuGet-Paket **sqlite-net-pcl**, wählen Sie es aus, und klicken Sie auf **Paket hinzufügen**, um es dem Projekt hinzuzufügen:

    ![](database-images/vsmac/add-package.png "Paket hinzufügen")

    > [!NOTE]
    > Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:
    > - **Autor:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Trotz des Paketnamens kann dieses NuGet-Paket in .NET Standard-Projekten verwendet werden.

    Mit diesem Paket können Datenbankvorgänge in der Anwendung verwendet werden.

4. Öffnen Sie im **Lösungspad**im Projekt " **Notizen** " **Note.cs** im Ordner " **Models** ", und ersetzen Sie den vorhandenen Code durch den folgenden Code:

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

    Diese Klasse definiert ein `Note` Modell, das Daten zu jeder Notiz in der Anwendung speichert. Die `ID` -Eigenschaft ist mit `PrimaryKey` dem `AutoIncrement` -Attribut und dem- `Note` Attribut gekennzeichnet, um sicherzustellen, dass jede Instanz in der SQLite.net-Datenbank eine eindeutige ID hat, die von SQLite.net.

    Speichern Sie die Änderungen an **Note.cs**, indem Sie **Datei > Speichern** auswählen (oder indem Sie  **&#8984; + S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

5. Fügen Sie im **Lösungspad** dem **Notes** -Projekt einen neuen Ordner mit dem Namen " **Data** " hinzu.

6. Fügen Sie im **Lösungspad** im Projekt **Notes** dem Ordner **Data** eine neue Klasse mit dem Namen **notedatabase** hinzu.

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

    Diese Klasse enthält Code zum Erstellen der Datenbank, zum Lesen von Daten, zum Schreiben von Daten in die Datenbank und zum Löschen von Daten aus der Datenbank. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `NoteDatabase`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad wird von der `App` -Klasse im nächsten Schritt bereitgestellt.

    Speichern Sie die Änderungen an **NoteDatabase.cs**, indem Sie **Datei > Speichern** auswählen (oder indem Sie  **&#8984; + S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

8. Doppelklicken Sie im **Lösungspad** im Projekt **Notes** auf **app.XAML.cs**, um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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

    Dieser Code definiert eine `Database` Eigenschaft, die eine neue `NoteDatabase` Instanz als Singleton erstellt, wobei der Dateiname der Datenbank als Argument an den `NoteDatabase` Konstruktor übergeben wird. Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

9. Doppelklicken Sie im **Lösungspad** im Projekt **Notes** auf **NotesPage.XAML.cs**, um es zu öffnen. Ersetzen Sie anschließend `OnAppearing` die-Methode durch den folgenden Code:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Dieser Code füllt den [`ListView`](xref:Xamarin.Forms.ListView) mit allen in der Datenbank gespeicherten Notizen auf.

    Speichern Sie die Änderungen an **NotesPage.XAML.cs** , indem Sie **Datei > Speichern** auswählen (oder indem Sie  **&#8984; + S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

10. Doppelklicken Sie im **Lösungspad** auf **NoteEntryPage.XAML.cs**, um es zu öffnen. Ersetzen Sie anschließend `OnSaveButtonClicked` die `OnDeleteButtonClicked` -Methode und die-Methode durch den folgenden Code:

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

      Speichert eine `Note` -Instanz, die einen einzelnen Hinweis darstellt, in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite. `NoteEntryPage` Wenn der `OnSaveButtonClicked` Ereignishandler ausgeführt wird, wird `Note` die Instanz in der Datenbank gespeichert, und die Anwendung navigiert zurück zur vorherigen Seite. Wenn der `OnDeleteButtonClicked` Ereignishandler ausgeführt wird, wird `Note` die Instanz aus der Datenbank gelöscht, und die Anwendung navigiert zurück zur vorherigen Seite.

      Speichern Sie die Änderungen an **NoteEntryPage.XAML.cs**, indem Sie **Datei > Speichern** auswählen (oder indem Sie  **&#8984; + S** drücken), und schließen Sie die Datei.

11. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter Erste Schritte mit [der Schnellstartanleitung](single-page.md#building-the-quickstart).

    Klicken Sie auf der **Seite NotesPage** auf die **+** Schaltfläche, um zur **noteentrypage** zu navigieren, und geben Sie einen Hinweis ein. Nach dem Speichern des Hinweises wird die Anwendung zurück zur **NotesPage** navigiert.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes gelernt:

- Verwenden Sie den nuget-Paket-Manager, um ein nuget-Paket zu einem Projekt hinzuzufügen.
- Lokale Speicherung von Daten in einer SQLite.net-Datenbank.

Wenn Sie die Anwendung mit XAML-Stilen formatieren möchten, fahren Sie mit der nächsten Schnellstartanleitung fort.

> [!div class="nextstepaction"]
> [Nächste](styling.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Xamarin. Forms-Schnellstart Deep Dive](deepdive.md)
