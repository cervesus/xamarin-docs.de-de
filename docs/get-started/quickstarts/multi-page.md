---
title: Navigieren in einer Xamarin.Forms-Multi-Page-Anwendung
description: In diesem Artikel wird erläutert, wie Sie die Single-Page-Anwendung, die nur eine einzige Notiz speichern kann, in eine Multi-Page-Anwendung umwandeln können, in der mehrere Notizen gespeichert werden können.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8858a66d2caeea815bf06d5d2741bfc3623598f5
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933730"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>Navigieren in einer Xamarin.Forms-Multi-Page-Anwendung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

In diesem Schnellstart lernen Sie Folgendes:

- Hinzufügen weiterer Seiten zu einer Xamarin.Forms-Projektmappe
- Navigieren zwischen Seiten
- Verwenden von Datenbindungen zum Synchronisieren von Daten zwischen Benutzeroberflächenelementen und ihren Datenquellen

In diesem Schnellstart erfahren Sie, wie Sie eine plattformübergreifende Xamarin.Forms-Single-Page-Anwendung, in der eine einzige Notiz gespeichert werden kann, in eine Multi-Page-Anwendung umwandeln, in der mehrere Notizen gespeichert werden können. Die fertige Anwendung wird unten gezeigt:

[![Notizenseite](multi-page-images/screenshots1-sml.png)](multi-page-images/screenshots1.png#lightbox "NotesPage")
[![Notizeneintragsseite](multi-page-images/screenshots2-sml.png)](multi-page-images/screenshots2.png#lightbox "NoteEntryPage")

### <a name="prerequisites"></a>Voraussetzungen

Sie sollten den [vorherigen Schnellstart](single-page.md) erfolgreich abgeschlossen haben, bevor Sie mit diesem Schnellstart beginnen. Alternativ können Sie auch das [letzte Schnellstartbeispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/) herunterladen und als Startpunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio. Klicken Sie im Startfenster in der Liste zuletzt verwendeter Projekte/Projektmappen auf die Projektmappe **Notizen**, oder klicken Sie auf die Option **Projekt oder Projektmappe öffnen**, und wählen Sie dann im Dialogfeld **Projekt oder Projektmappe öffnen** die Projektmappendatei für das Notizenprojekt aus:

    ![Projekt öffnen](multi-page-images/vs/open-solution.png)

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Notizen**, und klicken Sie dann auf **Hinzufügen > Neuer Ordner**:

    ![Neues Element hinzufügen](multi-page-images/vs/add-new-item.png)

3. Geben Sie im **Projektmappen-Explorer** dem neuen Ordner den Namen **Modelle**:

    ![Ordner „Modelle“](multi-page-images/vs/name-folder.png)

4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Modelle**, und klicken Sie anschließend auf **Hinzufügen > Neues Element**:

    ![Neue Datei hinzufügen](multi-page-images/vs/add-new-models-file.png)

5. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Elemente > Klasse**, nennen Sie die neue Datei **Notizen**, und klicken Sie auf die Schaltfläche **Hinzufügen**:

    ![Klasse „Notiz“ hinzufügen](multi-page-images/vs/add-note-class.png)

    Dadurch wird im Ordner **Modelle** des Projekts **Notizen** eine Klasse namens **Notiz** hinzugefügt.

6. Entfernen Sie in der Datei **Note.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

    Diese Klasse definiert ein `Note`-Modell, das Daten über jede Notiz in der Anwendung speichert.    

    Speichern Sie die Änderungen an **Note.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

7. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Notizen**, und klicken Sie auf **Hinzufügen > Neues Element**: Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C#-Elemente > Xamarin.Forms > Inhaltsseite**, nennen Sie die neue Datei **NoteEntryPage**, und klicken Sie auf die Schaltfläche **Hinzufügen**:

    ![Hinzufügen eines Xamarin.Forms ContentPage-Elements](multi-page-images/vs/add-note-entry-page.png)

    Dadurch wird im Stammordner des Projekts eine neue Seite namens **NoteEntryPage** hinzugefügt. Diese Seite ist die zweite Seite in der Anwendung.

8. Entfernen Sie in **NoteEntryPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

      Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Editor`](xref:Xamarin.Forms.Editor)-Klasse für die Texteingabe und zwei [`Button`](xref:Xamarin.Forms.Button)-Instanzen besteht, die die Anwendung zum Speichern oder Löschen einer Datei anweisen. Die beiden `Button`-Instanzen sind horizontal in einer [`Grid`](xref:Xamarin.Forms.Grid)-Klasse angeordnet, wobei `Editor` und `Grid` vertikal in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse angeordnet sind. Außerdem verwendet `Editor` Datenbindung, um die `Text`-Eigenschaft des `Note`-Modells zu binden. Weitere Informationen zur Datenbindung finden Sie im Abschnitt [Datenbindung](deepdive.md#data-binding) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

9. Entfernen Sie dann in **NoteEntryPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

      Dieser Code speichert in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite eine `Note`-Instanz, die für eine einzelne Notiz steht. Wenn auf die [`Button`](xref:Xamarin.Forms.Button) zum **Speichern** geklickt wird, wird der `OnSaveButtonClicked`-Ereignishandler ausgeführt. Entweder wird dann der `Editor`-Inhalt in eine neue Datei unter einem zufällig generierten Dateinamen gespeichert, oder in eine vorhandene Datei, wenn eine Notiz aktualisiert wird. In beiden Fällen wird die Datei im Ordner mit lokalen Anwendungsdaten der Anwendung gespeichert. Dann navigiert die Methode zurück zur vorherigen Seite. Wenn auf die **Löschen**-`Button` geklickt wird, wird der `OnDeleteButtonClicked`-Ereignishandler ausgeführt, sodass die Datei (falls vorhanden) gelöscht wird. Anschließend erfolgt die Navigation zurück zur vorherigen Seite. Weitere Informationen zur Navigation finden Sie im Abschnitt [Navigation](deepdive.md#navigation) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

      > [!WARNING]
      > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

10. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Notizen**, und klicken Sie auf **Hinzufügen > Neues Element**: Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C#-Elemente > Xamarin.Forms > Inhaltsseite**, nennen Sie die neue Datei **NotesPage**, und klicken Sie auf die Schaltfläche **Hinzufügen**.

      Dadurch wird im Stammordner des Projekts eine Seite namens **NotesPage** hinzugefügt. Diese Seite ist die Stammseite der Anwendung.

11. Entfernen Sie in **NotesPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die aus den Klassen [`ListView`](xref:Xamarin.Forms.ListView) und [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) besteht. `ListView` verwendet Datenbindung, um Notizen anzuzeigen, die von der Anwendung abgerufen werden. Das Auswählen einer Notiz führt zur Navigation zu `NoteEntryPage`. Dort kann die Notiz bearbeitet werden. Alternativ kann auch eine neue Notiz erstellt werden, indem Sie auf `ToolbarItem` klicken. Weitere Informationen zur Datenbindung finden Sie im Abschnitt [Datenbindung](deepdive.md#data-binding) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

12. Entfernen Sie in **NotesPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

    Dieser Code definiert die Funktionalität für `NotesPage`. Wenn die Seite angezeigt wird, wird die `OnAppearing`-Methode ausgeführt, die in [`ListView`](xref:Xamarin.Forms.ListView) alle Notizen lädt, die aus dem Ordner mit lokalen Anwendungsdaten abgerufen wurden. Wenn auf die [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)-Klasse geklickt wird, wird der `OnNoteAddedClicked`-Ereignishandler ausgeführt. Diese Methode navigiert zu `NoteEntryPage`. Dabei wird die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft von `NoteEntryPage` auf eine neue `Note`-Instanz festgelegt. Wenn ein Element von `ListView` ausgewählt wird, wird der `OnListViewItemSelected`-Ereignishandler ausgeführt. Diese Methode navigiert zu `NoteEntryPage`. Dabei wird die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft von `NoteEntryPage` auf die ausgewählte `Note`-Instanz festgelegt. Weitere Informationen zur Navigation finden Sie im Abschnitt [Navigation](deepdive.md#navigation) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

13. Doppelklicken Sie im **Projektmappen-Explorer** auf **App.xaml.cs**, um die Datei zu öffnen. Ersetzen Sie dann den vorhandenen Code durch folgenden Code:

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

    Dieser Code fügt eine Namespacedeklaration für den `System.IO`-Namespace hinzu, und fügt eine Deklaration für eine statische `FolderPath`-Eigenschaft mit dem Typ `string` hinzu. Die `FolderPath`-Eigenschaft wird verwendet, um den Pfad auf dem Gerät zum Speicherort zu speichern, an dem die Notizendaten gespeichert werden. Außerdem initialisiert der Code die `FolderPath`-Eigenschaft im `App`-Konstruktor und initialisiert die [`MainPage`](xref:Xamarin.Forms.Application.MainPage)-Eigenschaft als [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse, auf der eine Instanz von `NotesPage` gehostet wird. Weitere Informationen zur Navigation finden Sie im Abschnitt [Navigation](deepdive.md#navigation) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

14. Klicken Sie im **Projektmappen-Explorer** im Projekt **Notizen** mit der rechten Maustaste auf die Datei **MainPage.xaml**, und klicken Sie dann auf **Löschen**. Klicken Sie im Dialogfeld, das angezeigt wird, auf die Schaltfläche **OK**, damit die Datei von Ihrer Festplatte entfernt wird.

    Dadurch wird eine Seite entfernt, die nicht mehr verwendet wird.

15. Erstellen Sie das Projekt, und führen Sie es für jede Plattform aus. Weitere Informationen finden Sie unter [Erstellen des Schnellstarts](single-page.md#building-the-quickstart).

    Klicken Sie auf der **NotesPage**-Eigenschaft auf die Schaltfläche **+** , um zur Eigenschaft **NoteEntryPage** zu navigieren und eine Notiz einzugeben. Nach dem Speichern der Notiz navigiert die Anwendung zurück zur Eigenschaft **NotesPage**.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac. Klicken Sie auf der Startseite auf **Öffnen**, und wählen Sie im Dialogfeld die Projektmappendatei für das Notizenprojekt aus:

    ![Projektmappe öffnen](multi-page-images/vsmac/open-solution.png)

2. Wählen Sie im **Lösungspad** das Projekt **Notes** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neuer Ordner**:

    ![Neuen Ordner hinzufügen](multi-page-images/vsmac/add-new-folder.png)

3. Geben Sie im **Lösungspad** dem neuen Ordner den Namen **Modelle**:

    ![Ordner „Modelle“](multi-page-images/vsmac/name-folder.png)

4. Wählen Sie im **Lösungspad** das Projekt **Modelle** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…** :

    ![Neue Datei hinzufügen](multi-page-images/vsmac/add-new-models-file.png)

5. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **Notiz**, und klicken Sie auf die Schaltfläche **Neu**:

    ![Klasse „Notiz“ hinzufügen](multi-page-images/vsmac/add-note-class.png)

    Dadurch wird im Ordner **Modelle** des Projekts **Notizen** eine Klasse namens **Notiz** hinzugefügt.

6. Entfernen Sie in der Datei **Note.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

    Diese Klasse definiert ein `Note`-Modell, das Daten über jede Notiz in der Anwendung speichert.

    Speichern Sie die Änderungen an **Note.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

7. Wählen Sie im **Lösungspad** das Projekt **Notizen** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…** . Klicken Sie im Dialogfeld **Neue Datei** auf **Formular > XAML für Formular-ContentPage**, nennen Sie die neue Datei **NoteEntryPage**, und klicken Sie auf die Schaltfläche **Neu**:

    ![Hinzufügen eines Xamarin.Forms ContentPage-Elements](multi-page-images/vsmac/add-note-entry-page.png)

    Dadurch wird im Stammordner des Projekts eine neue Seite namens **NoteEntryPage** hinzugefügt. Diese Seite ist die zweite Seite in der Anwendung.

8. Entfernen Sie in **NoteEntryPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

      Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Editor`](xref:Xamarin.Forms.Editor)-Klasse für die Texteingabe und zwei [`Button`](xref:Xamarin.Forms.Button)-Instanzen besteht, die die Anwendung zum Speichern oder Löschen einer Datei anweisen. Die beiden `Button`-Instanzen sind horizontal in einer [`Grid`](xref:Xamarin.Forms.Grid)-Klasse angeordnet, wobei `Editor` und `Grid` vertikal in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse angeordnet sind. Außerdem verwendet `Editor` Datenbindung, um die `Text`-Eigenschaft des `Note`-Modells zu binden. Weitere Informationen zur Datenbindung finden Sie im Abschnitt [Datenbindung](deepdive.md#data-binding) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

9. Entfernen Sie dann in **NoteEntryPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

      Dieser Code speichert in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite eine `Note`-Instanz, die für eine einzelne Notiz steht. Wenn auf die [`Button`](xref:Xamarin.Forms.Button) zum **Speichern** geklickt wird, wird der `OnSaveButtonClicked`-Ereignishandler ausgeführt. Entweder wird dann der `Editor`-Inhalt in eine neue Datei unter einem zufällig generierten Dateinamen gespeichert, oder in eine vorhandene Datei, wenn eine Notiz aktualisiert wird. In beiden Fällen wird die Datei im Ordner mit lokalen Anwendungsdaten der Anwendung gespeichert. Dann navigiert die Methode zurück zur vorherigen Seite. Wenn auf die **Löschen**-`Button` geklickt wird, wird der `OnDeleteButtonClicked`-Ereignishandler ausgeführt, sodass die Datei (falls vorhanden) gelöscht wird. Anschließend erfolgt die Navigation zurück zur vorherigen Seite. Weitere Informationen zur Navigation finden Sie im Abschnitt [Navigation](deepdive.md#navigation) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

      > [!WARNING]
      > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

10. Wählen Sie im **Lösungspad** das Projekt **Notizen** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…** . Klicken Sie im Dialogfeld **Neue Datei** auf **Formular > XAML für Formular-ContentPage**, nennen Sie die neue Datei **NotesPage**, und klicken Sie auf die Schaltfläche **Neu**.

      Dadurch wird im Stammordner des Projekts eine Seite namens **NotesPage** hinzugefügt. Diese Seite ist die Stammseite der Anwendung.

11. Entfernen Sie in **NotesPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die aus den Klassen [`ListView`](xref:Xamarin.Forms.ListView) und [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) besteht. `ListView` verwendet Datenbindung, um Notizen anzuzeigen, die von der Anwendung abgerufen werden. Das Auswählen einer Notiz führt zur Navigation zu `NoteEntryPage`. Dort kann die Notiz bearbeitet werden. Alternativ kann auch eine neue Notiz erstellt werden, indem Sie auf `ToolbarItem` klicken. Weitere Informationen zur Datenbindung finden Sie im Abschnitt [Datenbindung](deepdive.md#data-binding) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

12. Entfernen Sie in **NotesPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

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

    Dieser Code definiert die Funktionalität für `NotesPage`. Wenn die Seite angezeigt wird, wird die `OnAppearing`-Methode ausgeführt, die in [`ListView`](xref:Xamarin.Forms.ListView) alle Notizen lädt, die aus dem Ordner mit lokalen Anwendungsdaten abgerufen wurden. Wenn auf die [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)-Klasse geklickt wird, wird der `OnNoteAddedClicked`-Ereignishandler ausgeführt. Diese Methode navigiert zu `NoteEntryPage`. Dabei wird die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft von `NoteEntryPage` auf eine neue `Note`-Instanz festgelegt. Wenn ein Element von `ListView` ausgewählt wird, wird der `OnListViewItemSelected`-Ereignishandler ausgeführt. Diese Methode navigiert zu `NoteEntryPage`. Dabei wird die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft von `NoteEntryPage` auf die ausgewählte `Note`-Instanz festgelegt. Weitere Informationen zur Navigation finden Sie im Abschnitt [Navigation](deepdive.md#navigation) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!WARNING]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die in weiteren Schritten behoben werden.

13. Doppelklicken Sie im **Lösungspad** auf **App.xaml.cs**, um die Datei zu öffnen. Ersetzen Sie dann den vorhandenen Code durch folgenden Code:

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

    Dieser Code fügt eine Namespacedeklaration für den `System.IO`-Namespace hinzu, und fügt eine Deklaration für eine statische `FolderPath`-Eigenschaft mit dem Typ `string` hinzu. Die `FolderPath`-Eigenschaft wird verwendet, um den Pfad auf dem Gerät zum Speicherort zu speichern, an dem die Notizendaten gespeichert werden. Außerdem initialisiert der Code die `FolderPath`-Eigenschaft im `App`-Konstruktor und initialisiert die [`MainPage`](xref:Xamarin.Forms.Application.MainPage)-Eigenschaft als [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse, auf der eine Instanz von `NotesPage` gehostet wird. Weitere Informationen zur Navigation finden Sie im Abschnitt [Navigation](deepdive.md#navigation) des Artikels [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

14. Klicken Sie im **Projektmappen-Explorer** im Projekt **Notizen** mit der rechten Maustaste auf die Datei **MainPage.xaml**, und klicken Sie dann auf **Entfernen**. Klicken Sie im Dialogfeld, das angezeigt wird, auf die Schaltfläche **Löschen**, damit die Datei von Ihrer Festplatte entfernt wird.

    Dadurch wird eine Seite entfernt, die nicht mehr verwendet wird.

15. Erstellen Sie das Projekt, und führen Sie es für jede Plattform aus. Weitere Informationen finden Sie unter [Erstellen des Schnellstarts](single-page.md#building-the-quickstart).

    Klicken Sie auf der **NotesPage**-Eigenschaft auf die Schaltfläche **+** , um zur Eigenschaft **NoteEntryPage** zu navigieren und eine Notiz einzugeben. Nach dem Speichern der Notiz navigiert die Anwendung zurück zur Eigenschaft **NotesPage**.

    Geben Sie eine Anzahl von Notizen unterschiedlicher Länge ein, um das Verhalten der Anwendung zu beobachten.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

- Hinzufügen weiterer Seiten zu einer Xamarin.Forms-Projektmappe
- Navigieren zwischen Seiten
- Verwenden von Datenbindungen zum Synchronisieren von Daten zwischen Benutzeroberflächenelementen und ihren Datenquellen

Wenn Sie die Anwendung so bearbeiten möchten, dass ihre Daten in einer lokalen SQLite.NET-Datenbank gespeichert werden, fahren Sie mit dem nächsten Schnellstart fort.

> [!div class="nextstepaction"]
> [Nächste](database.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md)
