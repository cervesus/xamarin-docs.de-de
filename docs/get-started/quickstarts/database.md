---
title: Speichern von Daten in einer lokalen SQLite.NET-Datenbank
description: In diesem Artikel wird erläutert, wie Daten in einer lokalen von SQLite.NET-Datenbank gespeichert.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: ebf0f21ed57b7d436721018abb2dca329b56baa4
ms.sourcegitcommit: 215b507b2e5a44bb023abc2c804c824b1a6190d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67194959"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>Daten in einer lokalen Datenbank für die von SQLite.NET Store

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/)

In dieser schnellstartanleitung erfahren Sie, wie Sie:

- Verwenden Sie den NuGet-Paket-Manager, um ein NuGet-Paket zu einem Projekt hinzuzufügen.
- Store Sie Daten lokal in einer Datenbank von SQLite.NET.

Der Schnellstart erläutert, wie Daten in einer lokalen von SQLite.NET-Datenbank gespeichert. Die fertige Anwendung wird unten gezeigt:

[![](database-images/screenshots1-sml.png "Anmerkungen zu dieser Seite")](database-images/screenshots1.png#lightbox "– Anmerkungen zu dieser Seite")
[![](database-images/screenshots2-sml.png "Beachten Sie die Seite für")](database-images/screenshots2.png#lightbox "Hinweis Seite für")

### <a name="prerequisites"></a>Vorraussetzungen

Sie sollte erfolgreich abgeschlossen. die [vorherigen schnellstartanleitung](multi-page.md) bevor Sie versuchen, die in dieser schnellstartanleitung. Alternativ Herunterladen der [vorherigen schnellstartbeispiel](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/) und als Ausgangspunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio, und öffnen Sie die Anmerkungen zu dieser Lösung.

2. In **Projektmappen-Explorer**, wählen die **Anmerkungen zu dieser** -Projekts mit der rechten Maustaste, und wählen **NuGet-Pakete verwalten...** :

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

4. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** -Projekt, öffnen **Note.cs** in die **Modelle** Ordner, und Ersetzen Sie den vorhandenen code, mit der folgender Code:

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

    Diese Klasse definiert eine `Note` Modell, das Daten über jeden Hinweis in der Anwendung gespeichert werden. Die `ID` Eigenschaft ist mit markiert `PrimaryKey` und `AutoIncrement` Attribute sicher, dass `Note` Instanz in der Datenbank von SQLite.NET wird eine eindeutige Id, die von von SQLite.NET bereitgestellt haben.

    Speichern Sie die Änderungen an **Note.cs** durch Drücken von **STRG + S**, und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

5. In **Projektmappen-Explorer**, fügen Sie einen neuen Ordner namens **Daten** auf die **Anmerkungen zu dieser** Projekt.

6. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** -Projekt fügen Sie eine neue Klasse mit dem Namen **NoteDatabase** auf die **Daten** Ordner.

7. In **NoteDatabase.cs**, ersetzen Sie den vorhandenen Code durch den folgenden Code:

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

    Diese Klasse enthält Code, um die Datenbank erstellen, Daten daraus zu lesen, Schreiben von Daten, und Daten daraus zu löschen. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `NoteDatabase`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad erfolgt durch die `App` Klasse im nächsten Schritt.

    Speichern Sie die Änderungen an **NoteDatabase.cs** durch Drücken von **STRG + S**, und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

8. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **"App.Xaml.cs"** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
            //...
        }
    }
    ```

    Dieser Code definiert eine `Database` -Eigenschaft, die eine neue erstellt `NoteDatabase` Instanz als Singleton, übergeben den Dateinamen der Datenbank als Argument für die `NoteDatabase` Konstruktor. Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

9. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **NotesPage.xaml.cs** um ihn zu öffnen. Ersetzen Sie dann die `OnAppearing` Methode durch den folgenden Code:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Dieser Code füllt die [ `ListView` ](xref:Xamarin.Forms.ListView) mit Anmerkungen in der Datenbank gespeichert.

    Speichern Sie die Änderungen an **NotesPage.xaml.cs** durch Drücken von **STRG + S**, und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

10. In **Projektmappen-Explorer**, doppelklicken Sie auf **NoteEntryPage.xaml.cs** um ihn zu öffnen. Ersetzen Sie dann die `OnSaveButtonClicked` und `OnDeleteButtonClicked` Methoden mit den folgenden Code:

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

      Die `NoteEntryPage` speichert eine `Note` -Instanz, die in einer einzelnen Notiz, steht die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite. Wenn die `OnSaveButtonClicked` -Ereignishandler ausgeführt wird, die `Note` Instanz in der Datenbank gespeichert ist und die Anwendung navigiert zur vorherigen Seite zurück. Wenn die `OnDeleteButtonClicked` -Ereignishandler ausgeführt wird, die `Note` Instanz aus der Datenbank gelöscht, und die Anwendung navigiert zur vorherigen Seite zurück.

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs** durch Drücken von **STRG + S**, und schließen Sie die Datei.

11. Erstellen Sie und führen Sie das Projekt auf jeder Plattform. Weitere Informationen finden Sie unter [des Schnellstarts erstellen](single-page.md#building-the-quickstart).

    Auf der **NotesPage** drücken Sie die **+** Schaltfläche, um das Navigieren zu der **NoteEntryPage** und geben Sie einen Hinweis. Nach dem Speichern der Notiz. der Anwendungs navigiert zurück an die **NotesPage**.

    Geben Sie eine Anzahl von Anmerkungen zu dieser unterschiedlicher Länge, um das Verhalten der Anwendung zu beobachten.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und öffnen Sie das Notes-Projekt.

2. In der **Lösungspad**, wählen die **Anmerkungen zu dieser** -Projekts mit der rechten Maustaste, und wählen **hinzufügen > NuGet-Pakete hinzufügen...** :

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

4. In der **Lösungspad**in die **Anmerkungen zu dieser** -Projekt, öffnen **Note.cs** in die **Modelle** Ordner, und Ersetzen Sie den vorhandenen Code durch Folgendes Code:

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

    Diese Klasse definiert eine `Note` Modell, das Daten über jeden Hinweis in der Anwendung gespeichert werden. Die `ID` Eigenschaft ist mit markiert `PrimaryKey` und `AutoIncrement` Attribute sicher, dass `Note` Instanz in der Datenbank von SQLite.NET wird eine eindeutige Id, die von von SQLite.NET bereitgestellt haben.

    Speichern Sie die Änderungen an **Note.cs** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

5. In der **Lösungspad**, fügen Sie einen neuen Ordner namens **Daten** auf die **Anmerkungen zu dieser** Projekt.

6. In der **Lösungspad**in die **Anmerkungen zu dieser** -Projekt fügen Sie eine neue Klasse mit dem Namen **NoteDatabase** auf die **Daten** Ordner.

7. In **NoteDatabase.cs**, ersetzen Sie den vorhandenen Code durch den folgenden Code:

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

    Diese Klasse enthält Code, um die Datenbank erstellen, Daten daraus zu lesen, Schreiben von Daten, und Daten daraus zu löschen. Im Code werden asynchrone SQLite.NET-APIs verwendet, mit denen Datenbankvorgänge in Hintergrundthreads verschoben werden. Außerdem akzeptiert der `NoteDatabase`-Konstruktor den Pfad der Datenbankdatei als Argument. Dieser Pfad erfolgt durch die `App` Klasse im nächsten Schritt.

    Speichern Sie die Änderungen an **NoteDatabase.cs** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

8. In der **Lösungspad**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **"App.Xaml.cs"** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
            ...
        }
    }
    ```

    Dieser Code definiert eine `Database` -Eigenschaft, die eine neue erstellt `NoteDatabase` Instanz als Singleton, übergeben den Dateinamen der Datenbank als Argument für die `NoteDatabase` Konstruktor. Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

9. In der **Lösungspad**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **NotesPage.xaml.cs** um ihn zu öffnen. Ersetzen Sie dann die `OnAppearing` Methode durch den folgenden Code:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Dieser Code füllt die [ `ListView` ](xref:Xamarin.Forms.ListView) mit Anmerkungen in der Datenbank gespeichert.

    Speichern Sie die Änderungen an **NotesPage.xaml.cs** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

10. In der **Lösungspad**, doppelklicken Sie auf **NoteEntryPage.xaml.cs** um ihn zu öffnen. Ersetzen Sie dann die `OnSaveButtonClicked` und `OnDeleteButtonClicked` Methoden mit den folgenden Code:

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

      Die `NoteEntryPage` speichert eine `Note` -Instanz, die in einer einzelnen Notiz, steht die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite. Wenn die `OnSaveButtonClicked` -Ereignishandler ausgeführt wird, die `Note` Instanz in der Datenbank gespeichert ist und die Anwendung navigiert zur vorherigen Seite zurück. Wenn die `OnDeleteButtonClicked` -Ereignishandler ausgeführt wird, die `Note` Instanz aus der Datenbank gelöscht, und die Anwendung navigiert zur vorherigen Seite zurück.

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

11. Erstellen Sie und führen Sie das Projekt auf jeder Plattform. Weitere Informationen finden Sie unter [des Schnellstarts erstellen](single-page.md#building-the-quickstart).

    Auf der **NotesPage** drücken Sie die **+** Schaltfläche, um das Navigieren zu der **NoteEntryPage** und geben Sie einen Hinweis. Nach dem Speichern der Notiz. der Anwendungs navigiert zurück an die **NotesPage**.

    Geben Sie eine Anzahl von Anmerkungen zu dieser unterschiedlicher Länge, um das Verhalten der Anwendung zu beobachten.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In dieser schnellstartanleitung haben Sie gelernt, wie Sie:

- Verwenden Sie den NuGet-Paket-Manager, um ein NuGet-Paket zu einem Projekt hinzuzufügen.
- Store Sie Daten lokal in einer Datenbank von SQLite.NET.

Um die Anwendung mit XAML-Formatvorlagen zu formatieren, weiterhin mit dem nächsten Schnellstart fortfahren.

> [!div class="nextstepaction"]
> [Nächste](styling.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/)
- [Tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md)
