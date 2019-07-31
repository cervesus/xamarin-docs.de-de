---
title: Navigieren in einer Xamarin.Forms-App mit mehreren Seiten
description: In diesem Artikel wird erläutert, wie Sie die Single-Page-Anwendung, in der ein einzelner Hinweis gespeichert werden kann, in einer mehrseitigen Anwendung, in der mehrere Notizen gespeichert werden können, umwandeln können.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 9ce02b4c6412eab1f4b1003b262573c59940286c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653797"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>Ausführen der Navigation in einer mehrseitigen xamarin. Forms-Anwendung

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

In dieser Schnellstartanleitung erfahren Sie Folgendes:

- Fügen Sie einer xamarin. Forms-Projekt Mappe weitere Seiten hinzu.
- Führt die Navigation zwischen Seiten aus.
- Verwenden Sie die Datenbindung, um Daten zwischen Benutzeroberflächen Elementen und Ihrer Datenquelle zu synchronisieren.

In der Schnellstartanleitung erfahren Sie, wie Sie eine plattformübergreifende xamarin. Forms-Anwendung mit einer einzelnen Seite aktivieren, in der Sie einen einzelnen Hinweis in einer mehrseitigen Anwendung speichern können, um mehrere Notizen speichern zu können. Die fertige Anwendung wird unten gezeigt:

Seite "Notizen" [ ![(multi-page-images/screenshots1-sml.png " ")]] (multi-page-images/screenshots1.png#lightbox "Seite \"Notizen") " Notiz der Seite "Anmerkung der(multi-page-images/screenshots2.png#lightbox "") [Einstiegs ![(multi-page-images/screenshots2-sml.png " ")]Seite"] 


### <a name="prerequisites"></a>Vorraussetzungen

Sie sollten den [vorherigen Schnellstart](single-page.md) erfolgreich abgeschlossen haben, bevor Sie diesen Schnellstart durchführen. Alternativ können Sie das [vorherige Schnellstart Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/) herunterladen und als Ausgangspunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio. Klicken Sie im Startfenster in der Liste zuletzt verwendete Projekte/Projektmappen auf die Lösung **Notizen** , oder klicken Sie auf **Projekt oder Projekt Mappe öffnen**, und wählen Sie im Dialogfeld **Projekt/Projekt Mappe öffnen** die Projektmappendatei für das Notizen-Projekt aus:

    ![](multi-page-images/vs/open-solution.png "Projekt öffnen")

2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das **Notizen** -Projekt, und wählen Sie **> neuen Ordner hinzufügen**aus:

    ![](multi-page-images/vs/add-new-item.png "Neues Element hinzufügen")

3. Benennen Sie in **Projektmappen-Explorer**die neuen Ordner **Modelle**:

    ![](multi-page-images/vs/name-folder.png "Ordner \"Models\"")

4. Wählen Sie in **Projektmappen-Explorer**den Ordner **Modelle** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **> Neues Element hinzufügen...** aus:

    ![](multi-page-images/vs/add-new-models-file.png "Neue Datei hinzufügen")

5. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **visuelle C# Elemente > Klasse**aus, benennen Sie die **neue Datei,** und klicken Sie auf die Schaltfläche **Hinzufügen** :

    ![](multi-page-images/vs/add-note-class.png "Note-Klasse hinzufügen")

    Dadurch wird dem Ordner " **Models** " des **Notes** -Projekts eine Klasse mit dem Namen " **Note** " hinzugefügt.

6. Entfernen Sie in **Note.cs**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Diese Klasse definiert ein `Note` Modell, das Daten zu jeder Notiz in der Anwendung speichert.    

    Speichern Sie die Änderungen an **Note.cs** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

7. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das **Notizen** -Projekt, und wählen Sie **> Neues Element hinzufügen..** . aus. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **visuelle C# Elemente > xamarin. Forms > Seite Inhalt**aus, benennen Sie die neue Datei **noteentrypage**, und klicken Sie auf die Schaltfläche **Hinzufügen** :

    ![](multi-page-images/vs/add-note-entry-page.png "Xamarin. Forms-ContentPage hinzufügen")

    Dadurch wird dem Stamm Ordner des Projekts eine neue Seite mit dem Namen **noteentrypage** hinzugefügt. Diese Seite wird die zweite Seite in der Anwendung.

8. Entfernen Sie in **noteentrypage. XAML**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus [`Editor`](xref:Xamarin.Forms.Editor) für Texteingaben besteht, und zwei [`Button`](xref:Xamarin.Forms.Button) -Instanzen, die die Anwendung dazu leiten, eine Datei zu speichern oder zu löschen. Die beiden `Button` `Editor` -Instanzen werden horizontal in einem [`Grid`](xref:Xamarin.Forms.Grid)angeordnet, wobei und `Grid` vertikal in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout)angeordnet werden. Außerdem verwendet die `Editor` Datenbindung, um eine Bindung an die `Text` -Eigenschaft des `Note` Modells herzustellen. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding](deepdive.md#data-binding) in the [xamarin. Forms Quick Start Deep Dive](deepdive.md).

      Speichern Sie die Änderungen an **noteentrypage. XAML** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

9. Entfernen Sie in **NoteEntryPage.XAML.cs**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      In diesem Code wird `Note` eine-Instanz, die einen einzelnen Hinweis darstellt, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) in der der Seite gespeichert. Beim Drücken der **Speicherung** [`Button`](xref:Xamarin.Forms.Button) wird der `OnSaveButtonClicked` -Ereignishandler ausgeführt, bei `Editor` dem der Inhalt der in einer neuen Datei mit einem zufällig generierten Dateinamen gespeichert wird, oder in einer vorhandenen Datei, wenn ein Hinweis aktualisiert wird. In beiden Fällen wird die Datei im Ordner Lokale Anwendungsdaten für die Anwendung gespeichert. Anschließend navigiert die Methode zurück zur vorherigen Seite. Beim `Button` drücken **des Lösch** Vorgängen wird der Ereignishandlerausgeführt,derdieDateilöscht(sofernvorhanden)undzurvorherigenSeitezurücknavigiert.`OnDeleteButtonClicked` Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in [xamarin. Forms Quick Start Deep Dive](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.XAML.cs** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

      > [!WARNING]
      > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

10. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das **Notizen** -Projekt, und wählen Sie **> Neues Element hinzufügen..** . aus. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **visuelle C# Elemente > xamarin. Forms > Seite Inhalt**aus, benennen Sie die neue Datei in **NotesPage**, und klicken Sie auf die Schaltfläche **Hinzufügen** .

      Dadurch wird dem Stamm Ordner des Projekts eine Seite mit dem Namen " **NotesPage** " hinzugefügt. Diese Seite ist die Stamm Seite der Anwendung.

11. Entfernen Sie in " **NotesPage. XAML**" den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Mit diesem Code wird die Benutzeroberfläche für die Seite deklarativ definiert, die aus [`ListView`](xref:Xamarin.Forms.ListView) einem und [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)einem besteht. Der `ListView` verwendet die Datenbindung, um alle von der Anwendung abgerufenen Notizen anzuzeigen. Wenn Sie eine Notiz auswählen, wird `NoteEntryPage` zu der navigiert, in der der Hinweis geändert werden kann. Alternativ kann ein neuer Hinweis durch Drücken `ToolbarItem`von erstellt werden. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding](deepdive.md#data-binding) in the [xamarin. Forms Quick Start Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an der Datei " **NotesPage. XAML** ", indem Sie **STRG + S**drücken, und schließen Sie die Datei.

12. Entfernen Sie in **NotesPage.XAML.cs**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Dieser Code definiert die Funktionalität für das `NotesPage`. Wenn die Seite angezeigt wird, `OnAppearing` wird die-Methode ausgeführt, die den [`ListView`](xref:Xamarin.Forms.ListView) mit allen Notizen auffüllt, die aus dem lokalen Anwendungsdatenordner abgerufen wurden. `OnNoteAddedClicked` Wenn gedrückt [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) wird, wird der-Ereignishandler ausgeführt. Diese Methode navigiert zum `NoteEntryPage`, wobei der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` auf eine neue `Note` -Instanz festgelegt wird. Wenn ein Element in der `ListView` ausgewählt wird, `OnListViewItemSelected` wird der Ereignishandler ausgeführt. Diese Methode navigiert zum `NoteEntryPage`, wobei der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` auf die ausgewählte `Note` Instanz festgelegt wird. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in [xamarin. Forms Quick Start Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.XAML.cs** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

13. Doppelklicken Sie in **Projektmappen-Explorer**auf **app.XAML.cs** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    Mit diesem Code wird eine Namespace Deklaration `System.IO` für den-Namespace hinzugefügt, und es `FolderPath` wird eine Deklaration für eine statische Eigenschaft des Typs `string`hinzugefügt. Die `FolderPath` -Eigenschaft wird zum Speichern des Pfads auf dem Gerät verwendet, auf dem Hinweis Daten gespeichert werden. Außerdem `FolderPath` initialisiert der Code die-Eigenschaft `App` im Konstruktor und initialisiert die [`MainPage`](xref:Xamarin.Forms.Application.MainPage) -Eigenschaft als eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , die eine Instanz von `NotesPage`hostet. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in [xamarin. Forms Quick Start Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

14. Klicken Sie in **Projektmappen-Explorer**im **Notes** -Projekt mit der rechten Maustaste auf **MainPage. XAML**, und wählen Sie **Löschen**aus. Klicken Sie im angezeigten Dialogfeld auf die Schaltfläche **OK** , um die Datei von der Festplatte zu entfernen.

    Dadurch wird eine Seite entfernt, die nicht mehr verwendet wird.

15. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter Erste Schritte mit [der Schnellstartanleitung](single-page.md#building-the-quickstart).

    Klicken Sie auf der **Seite NotesPage** auf die **+** Schaltfläche, um zur **noteentrypage** zu navigieren, und geben Sie einen Hinweis ein. Nach dem Speichern des Hinweises wird die Anwendung zurück zur **NotesPage**navigiert.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac. Klicken Sie im Startfenster auf **Öffnen**, und wählen Sie im Dialogfeld die Projektmappendatei für das Notes-Projekt aus:

    ![](multi-page-images/vsmac/open-solution.png "Projektmappe öffnen")

2. Wählen Sie im **Lösungspad**das Projekt **Notes** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **> neuen Ordner hinzufügen**aus:

    ![](multi-page-images/vsmac/add-new-folder.png "Neuen Ordner hinzufügen")

3. Benennen Sie im **Lösungspad**die neuen Ordner **Modelle**:

    ![](multi-page-images/vsmac/name-folder.png "Ordner \"Models\"")

4. Wählen Sie im **Lösungspad**den Ordner **Modelle** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **> neue Datei hinzufügen...** aus:

    ![](multi-page-images/vsmac/add-new-models-file.png "Neue Datei hinzufügen")

5. Klicken Sie im Dialogfeld **neue Datei** auf **Allgemein > leere Klasse**, benennen Sie die **neue Datei,** und klicken Sie auf die Schaltfläche **neu** :

    ![](multi-page-images/vsmac/add-note-class.png "Note-Klasse hinzufügen")

    Dadurch wird dem Ordner " **Models** " des **Notes** -Projekts eine Klasse mit dem Namen " **Note** " hinzugefügt.

6. Entfernen Sie in **Note.cs**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Diese Klasse definiert ein `Note` Modell, das Daten zu jeder Notiz in der Anwendung speichert.

    Speichern Sie die Änderungen an **Note.cs** , indem Sie **Datei > Speichern** auswählen (oder indem Sie  **&#8984; + S**drücken), und schließen Sie die Datei.

7. Wählen Sie im **Lösungspad**das Projekt **Notes** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **> neue Datei hinzufügen...** aus. Wählen Sie im Dialogfeld **neue Datei** die Option **Formulare > Forms ContentPage XAML**aus, benennen Sie die neue Datei **noteentrypage**, und klicken Sie auf die Schaltfläche **neu** :

    ![](multi-page-images/vsmac/add-note-entry-page.png "Xamarin. Forms-ContentPage hinzufügen")

    Dadurch wird dem Stamm Ordner des Projekts eine neue Seite mit dem Namen **noteentrypage** hinzugefügt. Diese Seite wird die zweite Seite in der Anwendung.

8. Entfernen Sie in **noteentrypage. XAML**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus [`Editor`](xref:Xamarin.Forms.Editor) für Texteingaben besteht, und zwei [`Button`](xref:Xamarin.Forms.Button) -Instanzen, die die Anwendung dazu leiten, eine Datei zu speichern oder zu löschen. Die beiden `Button` `Editor` -Instanzen werden horizontal in einem [`Grid`](xref:Xamarin.Forms.Grid)angeordnet, wobei und `Grid` vertikal in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout)angeordnet werden. Außerdem verwendet die `Editor` Datenbindung, um eine Bindung an die `Text` -Eigenschaft des `Note` Modells herzustellen. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding](deepdive.md#data-binding) in the [xamarin. Forms Quick Start Deep Dive](deepdive.md).

      Speichern Sie die Änderungen an **noteentrypage. XAML** , indem Sie **Datei > Speichern** (oder durch Drücken  **&#8984; von + S**) auswählen, und schließen Sie die Datei.

9. Entfernen Sie in **NoteEntryPage.XAML.cs**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      In diesem Code wird `Note` eine-Instanz, die einen einzelnen Hinweis darstellt, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) in der der Seite gespeichert. Beim Drücken der **Speicherung** [`Button`](xref:Xamarin.Forms.Button) wird der `OnSaveButtonClicked` -Ereignishandler ausgeführt, bei `Editor` dem der Inhalt der in einer neuen Datei mit einem zufällig generierten Dateinamen gespeichert wird, oder in einer vorhandenen Datei, wenn ein Hinweis aktualisiert wird. In beiden Fällen wird die Datei im Ordner Lokale Anwendungsdaten für die Anwendung gespeichert. Anschließend navigiert die Methode zurück zur vorherigen Seite. Beim `Button` drücken **des Lösch** Vorgängen wird der Ereignishandlerausgeführt,derdieDateilöscht(sofernvorhanden)undzurvorherigenSeitezurücknavigiert.`OnDeleteButtonClicked` Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in [xamarin. Forms Quick Start Deep Dive](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.XAML.cs** , indem Sie **Datei > Speichern** auswählen (oder indem Sie  **&#8984; + S**drücken), und schließen Sie die Datei.

      > [!WARNING]
      > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

10. Wählen Sie im **Lösungspad**das Projekt **Notes** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **> neue Datei hinzufügen...** aus. Wählen Sie im Dialogfeld **neue Datei** die Option **Formulare > Forms ContentPage XAML**aus, benennen Sie die neue Datei **NotesPage**, und klicken Sie auf die Schaltfläche **neu** .

      Dadurch wird dem Stamm Ordner des Projekts eine Seite mit dem Namen " **NotesPage** " hinzugefügt. Diese Seite ist die Stamm Seite der Anwendung.

11. Entfernen Sie in " **NotesPage. XAML**" den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Mit diesem Code wird die Benutzeroberfläche für die Seite deklarativ definiert, die aus [`ListView`](xref:Xamarin.Forms.ListView) einem und [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)einem besteht. Der `ListView` verwendet die Datenbindung, um alle von der Anwendung abgerufenen Notizen anzuzeigen. Wenn Sie eine Notiz auswählen, wird `NoteEntryPage` zu der navigiert, in der der Hinweis geändert werden kann. Alternativ kann ein neuer Hinweis durch Drücken `ToolbarItem`von erstellt werden. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding](deepdive.md#data-binding) in the [xamarin. Forms Quick Start Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an der Datei " **NotesPage. XAML** ", indem Sie **Datei > Speichern** (oder durch Drücken  **&#8984; von + S**) auswählen, und schließen Sie die Datei.

12. Entfernen Sie in **NotesPage.XAML.cs**den gesamten Vorlagen Code, und ersetzen Sie ihn durch den folgenden Code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Dieser Code definiert die Funktionalität für das `NotesPage`. Wenn die Seite angezeigt wird, `OnAppearing` wird die-Methode ausgeführt, die den [`ListView`](xref:Xamarin.Forms.ListView) mit allen Notizen auffüllt, die aus dem lokalen Anwendungsdatenordner abgerufen wurden. `OnNoteAddedClicked` Wenn gedrückt [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) wird, wird der-Ereignishandler ausgeführt. Diese Methode navigiert zum `NoteEntryPage`, wobei der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` auf eine neue `Note` -Instanz festgelegt wird. Wenn ein Element in der `ListView` ausgewählt wird, `OnListViewItemSelected` wird der Ereignishandler ausgeführt. Diese Methode navigiert zum `NoteEntryPage`, wobei der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` auf die ausgewählte `Note` Instanz festgelegt wird. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in [xamarin. Forms Quick Start Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.XAML.cs** , indem Sie **Datei > Speichern** auswählen (oder indem Sie  **&#8984; + S**drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in den nachfolgenden Schritten korrigiert werden.

13. Doppelklicken Sie im **Lösungspad**auf **app.XAML.cs** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    Mit diesem Code wird eine Namespace Deklaration `System.IO` für den-Namespace hinzugefügt, und es `FolderPath` wird eine Deklaration für eine statische Eigenschaft des Typs `string`hinzugefügt. Die `FolderPath` -Eigenschaft wird zum Speichern des Pfads auf dem Gerät verwendet, auf dem Hinweis Daten gespeichert werden. Außerdem `FolderPath` initialisiert der Code die-Eigenschaft `App` im Konstruktor und initialisiert die [`MainPage`](xref:Xamarin.Forms.Application.MainPage) -Eigenschaft als eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , die eine Instanz von `NotesPage`hostet. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in [xamarin. Forms Quick Start Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

14. Klicken Sie im **Lösungspad**im **Notes** -Projekt mit der rechten Maustaste auf **MainPage. XAML**, und wählen Sie **Entfernen**aus. Klicken Sie im angezeigten Dialogfeld auf die Schaltfläche **Löschen** , um die Datei von der Festplatte zu entfernen.

    Dadurch wird eine Seite entfernt, die nicht mehr verwendet wird.

15. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter Erste Schritte mit [der Schnellstartanleitung](single-page.md#building-the-quickstart).

    Klicken Sie auf der **Seite NotesPage** auf die **+** Schaltfläche, um zur **noteentrypage** zu navigieren, und geben Sie einen Hinweis ein. Nach dem Speichern des Hinweises wird die Anwendung zurück zur **NotesPage**navigiert.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes gelernt:

- Fügen Sie einer xamarin. Forms-Projekt Mappe weitere Seiten hinzu.
- Führt die Navigation zwischen Seiten aus.
- Verwenden Sie die Datenbindung, um Daten zwischen Benutzeroberflächen Elementen und Ihrer Datenquelle zu synchronisieren.

Wenn Sie die Anwendung so ändern möchten, dass Ihre Daten in einer lokalen sqlite.net-Datenbank gespeichert werden, fahren Sie mit der nächsten Schnellstartanleitung fort.

> [!div class="nextstepaction"]
> [Nächste](database.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Xamarin. Forms-Schnellstart Deep Dive](deepdive.md)
