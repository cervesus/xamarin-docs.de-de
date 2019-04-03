---
title: Navigieren in einer Xamarin.Forms-App mit mehreren Seiten
description: In diesem Artikel erläutert das Aktivieren der einseitigen Anwendung, kann das Speichern von einer einzelnen Notiz, in einer Anwendung auf mehreren Seiten, die mehrere Notizen speichern kann.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 855962560897789dadba535f69c4a7da42bb4742
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854976"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>Führen Sie die Navigation in einer mehrseitigen Xamarin.Forms-Anwendung

[![Downloadliste Beispiel](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)

In dieser schnellstartanleitung erfahren Sie, wie Sie:

- Fügen Sie zusätzliche Seiten zu einer Xamarin.Forms-Projektmappe hinzu.
- Führen Sie die Navigation zwischen Seiten.
- Verwenden Sie die Datenbindung zum Synchronisieren von Daten zwischen Elementen der Benutzeroberfläche und ihre Datenquelle ein.

Der Schnellstart erläutert, Informationen zum Aktivieren von einer einzelnen Seite plattformübergreifende Xamarin.Forms-Anwendung, Speichern einer einzelnen Notiz, in einer Anwendung auf mehreren Seiten, die mehrere Notizen speichern kann. Die fertige Anwendung wird unten gezeigt:

[![](multi-page-images/screenshots1-sml.png "Anmerkungen zu dieser Seite")](multi-page-images/screenshots1.png#lightbox "– Anmerkungen zu dieser Seite")
[![](multi-page-images/screenshots2-sml.png "Beachten Sie die Seite für")](multi-page-images/screenshots2.png#lightbox "Hinweis Seite für")

### <a name="prerequisites"></a>Vorraussetzungen

Sie sollte erfolgreich abgeschlossen. die [vorherigen schnellstartanleitung](single-page.md) bevor Sie versuchen, die in dieser schnellstartanleitung. Alternativ Herunterladen der [vorherigen schnellstartbeispiel](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/) und als Ausgangspunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio. Klicken Sie im Startfenster auf die **Anmerkungen zu dieser** -Lösung in der Liste der zuletzt verwendeten Projekte und Projektmappen, oder klicken Sie auf **öffnet ein Projekt oder Projektmappe**, und klicken Sie in der **Projekt/Projektmappe öffnen** Dialogfeld Wählen Sie die Projektmappendatei für das Projekt für die Anmerkungen zu dieser Version:

    ![](multi-page-images/vs/open-solution.png "Projekt öffnen")

2. In **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Anmerkungen zu dieser** Projekt, und wählen **hinzufügen > Neuer Ordner**:

    ![](multi-page-images/vs/add-new-item.png "Neues Element hinzufügen")

3. In **Projektmappen-Explorer**, nennen Sie diesen Ordner **Modelle**:

    ![](multi-page-images/vs/name-folder.png "Ordner \"Models\"")

4. In **Projektmappen-Explorer**, wählen die **Modelle** Ordner, die mit der rechten Maustaste und wählen Sie **hinzufügen > Neues Element...** :

    ![](multi-page-images/vs/add-new-models-file.png "Neue Datei hinzufügen")

5. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual C# Elemente >-Klasse**, nennen Sie die neue Datei **Hinweis**, und klicken Sie auf die **hinzufügen** Schaltfläche:

    ![](multi-page-images/vs/add-note-class.png "Hinweis hinzufügen")

    Dadurch wird eine Klasse namens hinzugefügt **Hinweis** auf die **Modelle** Ordner die **Anmerkungen zu dieser** Projekt.

6. In **Note.cs**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

    Diese Klasse definiert eine `Note` Modell, das Daten über jeden Hinweis in der Anwendung gespeichert werden.    

    Speichern Sie die Änderungen an **Note.cs** durch Drücken von **STRG + S**, und schließen Sie die Datei.

7. In **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Anmerkungen zu dieser** Projekt, und wählen **hinzufügen > Neues Element...** . In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual C# Elemente > Xamarin.Forms > Inhaltsseite**, nennen Sie die neue Datei **NoteEntryPage**, und klicken Sie auf die  **Hinzufügen** Schaltfläche:

    ![](multi-page-images/vs/add-note-entry-page.png "Xamarin.Forms-ContentPage hinzufügen")

    Dadurch wird eine neue Seite mit dem Namen hinzugefügt **NoteEntryPage** in den Stammordner des Projekts. Auf dieser Seite werden der zweiten Seite in der Anwendung.

8. In **NoteEntryPage.xaml**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

      Dieser Code definiert deklarativ die Benutzeroberfläche auf der Seite besteht aus einem [ `Editor` ](xref:Xamarin.Forms.Editor) für Texteingabe und die beiden [ `Button` ](xref:Xamarin.Forms.Button) Instanzen, die die Anwendung zu speichern oder Löschen von weiterleiten eine Datei. Die beiden `Button` Instanzen horizontal angeordnet wird einem [ `Grid` ](xref:Xamarin.Forms.Grid), mit der `Editor` und `Grid` wird vertikal angeordnet, eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Darüber hinaus die `Editor` verwendet die Datenbindung zum Binden an die `Text` Eigenschaft der `Note` Modell. Weitere Informationen zur Datenbindung finden Sie unter [Datenbindung](deepdive.md#data-binding) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml** durch Drücken von **STRG + S**, und schließen Sie die Datei.

9. In **NoteEntryPage.xaml.cs**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

      Dieser Code speichert eine `Note` -Instanz, die in einer einzelnen Notiz, steht die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite. Wenn die **speichern** [ `Button` ](xref:Xamarin.Forms.Button) gedrückt wird die `OnSaveButtonClicked` -Ereignishandler ausgeführt wird, die entweder speichert den Inhalt des der `Editor` in eine neue Datei mit einem zufällig generierten Dateinamen, oder Um eine vorhandene Datei, wenn eine Notiz aktualisiert wird. In beiden Fällen wird die Datei im Ordner Data lokale Anwendung für die Anwendung gespeichert. Dann wird die Methode zurück zur vorherigen Seite navigiert. Wenn die **löschen** `Button` gedrückt wird die `OnDeleteButtonClicked` -Ereignishandler ausgeführt wird, das Sie die Datei, vorausgesetzt, dass es vorhanden ist, und Navigieren zurück zur vorherigen Seite gelöscht. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs** durch Drücken von **STRG + S**, und schließen Sie die Datei.

      > [!WARNING]
      > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

10. In **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Anmerkungen zu dieser** Projekt, und wählen **hinzufügen > Neues Element...** . In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual C# Elemente > Xamarin.Forms > Inhaltsseite**, nennen Sie die neue Datei **NotesPage**, und klicken Sie auf die **hinzufügen**  Schaltfläche.

      Dadurch wird eine Seite namens hinzugefügt **NotesPage** in den Stammordner des Projekts. Auf dieser Seite werden die Stammseite der Anwendung.

11. In **NotesPage.xaml**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

    Dieser Code definiert deklarativ die Benutzeroberfläche auf der Seite besteht aus einem [ `ListView` ](xref:Xamarin.Forms.ListView) und [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem). Die `ListView` verwendet Daten binden, um Hinweise anzuzeigen, die von der Anwendung abgerufen werden, und wählen eine Notiz werden, navigieren Sie zu der `NoteEntryPage` , in dem die Anmerkung kann geändert werden kann. Alternativ kann eine neue Notiz erstellt werden, durch Drücken der `ToolbarItem`. Weitere Informationen zur Datenbindung finden Sie unter [Datenbindung](deepdive.md#data-binding) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml** durch Drücken von **STRG + S**, und schließen Sie die Datei.

12. In **NotesPage.xaml.cs**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

    Dieser Code definiert die Funktionalität für die `NotesPage`. Wenn die Seite wird angezeigt, die `OnAppearing` Methode ausgeführt wird, wird die füllt die [ `ListView` ](xref:Xamarin.Forms.ListView) mit Anmerkungen, die aus dem lokalen Anwendungsordner für Daten abgerufen wurden. Wenn die [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) gedrückt wird die `OnNoteAddedClicked` -Ereignishandler ausgeführt wird. Diese Methode navigiert zu der `NoteEntryPage`wird durch das Festlegen der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von der `NoteEntryPage` in ein neues `Note` Instanz. Wenn ein Element in der `ListView` ausgewählt ist die `OnListViewItemSelected` -Ereignishandler ausgeführt wird. Diese Methode navigiert zu der `NoteEntryPage`wird durch das Festlegen der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von der `NoteEntryPage` , die dem ausgewählten `Note` Instanz. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml.cs** durch Drücken von **STRG + S**, und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

13. In **Projektmappen-Explorer**, doppelklicken Sie auf **"App.Xaml.cs"** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
            ...
        }
    }
    ```

    Dieser Code Fügt eine Namespacedeklaration für die `System.IO` -Namespace, und fügt eine Deklaration für eine statische `FolderPath` Eigenschaft vom Typ `string`. Die `FolderPath` Eigenschaft wird verwendet, um den Pfad auf dem Gerät gespeichert, in denen sich Daten gespeichert werden. Darüber hinaus, dass der Code initialisiert die `FolderPath` -Eigenschaft in der `App` Konstruktor und initialisiert die [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) Eigenschaft eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) , hostet ein Instanz von `NotesPage`. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

14. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** Projekt, mit der rechten Maustaste **"MainPage.xaml"**, und wählen Sie **löschen**. In der drücken Sie die angezeigten Dialogfeld die **OK** Schaltfläche, um die Datei von der Festplatte zu entfernen.

    Dadurch wird eine Seite, die nicht mehr verwendet wird, entfernt.

15. Erstellen Sie und führen Sie das Projekt auf jeder Plattform. Weitere Informationen finden Sie unter [des Schnellstarts erstellen](single-page.md#building-the-quickstart).

    Auf der **NotesPage** drücken Sie die **+** Schaltfläche, um das Navigieren zu der **NoteEntryPage** und geben Sie einen Hinweis. Nach dem Speichern der Notiz. der Anwendungs navigiert zurück an die **NotesPage**.

    Geben Sie eine Anzahl von Anmerkungen zu dieser unterschiedlicher Länge, um das Verhalten der Anwendung zu beobachten.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac. Klicken Sie im Startfenster auf **öffnen**, und wählen Sie im Dialogfeld die Projektmappendatei für das Projekt für die Anmerkungen zu dieser Version:

    ![](multi-page-images/vsmac/open-solution.png "Projektmappe öffnen")

2. In der **Lösungspad**, wählen die **Anmerkungen zu dieser** -Projekt, mit der rechten Maustaste und wählen Sie **hinzufügen > Neuer Ordner**:

    ![](multi-page-images/vsmac/add-new-folder.png "Hinzufügen eines neuen Ordners")

3. In der **Lösungspad**, nennen Sie diesen Ordner **Modelle**:

    ![](multi-page-images/vsmac/name-folder.png "Ordner \"Models\"")

4. In der **Lösungspad**, wählen die **Modelle** Ordner, die mit der rechten Maustaste und wählen Sie **hinzufügen > neue Datei...** :

    ![](multi-page-images/vsmac/add-new-models-file.png "Neue Datei hinzufügen")

5. In der **neue Datei** wählen Sie im Dialogfeld **Allgemein > leere Klasse**, nennen Sie die neue Datei **Hinweis**, und klicken Sie auf die **neu** Schaltfläche:

    ![](multi-page-images/vsmac/add-note-class.png "Hinweis hinzufügen")

    Dadurch wird eine Klasse namens hinzugefügt **Hinweis** auf die **Modelle** Ordner die **Anmerkungen zu dieser** Projekt.

6. In **Note.cs**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

    Diese Klasse definiert eine `Note` Modell, das Daten über jeden Hinweis in der Anwendung gespeichert werden.

    Speichern Sie die Änderungen an **Note.cs** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

7. In der **Lösungspad**, wählen die **Anmerkungen zu dieser** -Projekt, mit der rechten Maustaste und wählen Sie **hinzufügen > neue Datei...** . In der **neue Datei** wählen Sie im Dialogfeld **Forms > Forms-ContentPage XAML**, nennen Sie die neue Datei **NoteEntryPage**, und klicken Sie auf die **neu** Schaltfläche:

    ![](multi-page-images/vsmac/add-note-entry-page.png "Xamarin.Forms-ContentPage hinzufügen")

    Dadurch wird eine neue Seite mit dem Namen hinzugefügt **NoteEntryPage** in den Stammordner des Projekts. Auf dieser Seite werden der zweiten Seite in der Anwendung.

8. In **NoteEntryPage.xaml**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

      Dieser Code definiert deklarativ die Benutzeroberfläche auf der Seite besteht aus einem [ `Editor` ](xref:Xamarin.Forms.Editor) für Texteingabe und die beiden [ `Button` ](xref:Xamarin.Forms.Button) Instanzen, die die Anwendung zu speichern oder Löschen von weiterleiten eine Datei. Die beiden `Button` Instanzen horizontal angeordnet wird einem [ `Grid` ](xref:Xamarin.Forms.Grid), mit der `Editor` und `Grid` wird vertikal angeordnet, eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Darüber hinaus die `Editor` verwendet die Datenbindung zum Binden an die `Text` Eigenschaft der `Note` Modell. Weitere Informationen zur Datenbindung finden Sie unter [Datenbindung](deepdive.md#data-binding) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

9. In **NoteEntryPage.xaml.cs**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

      Dieser Code speichert eine `Note` -Instanz, die in einer einzelnen Notiz, steht die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite. Wenn die **speichern** [ `Button` ](xref:Xamarin.Forms.Button) gedrückt wird die `OnSaveButtonClicked` -Ereignishandler ausgeführt wird, die entweder speichert den Inhalt des der `Editor` in eine neue Datei mit einem zufällig generierten Dateinamen, oder Um eine vorhandene Datei, wenn eine Notiz aktualisiert wird. In beiden Fällen wird die Datei im Ordner Data lokale Anwendung für die Anwendung gespeichert. Dann wird die Methode zurück zur vorherigen Seite navigiert. Wenn die **löschen** `Button` gedrückt wird die `OnDeleteButtonClicked` -Ereignishandler ausgeführt wird, das Sie die Datei, vorausgesetzt, dass es vorhanden ist, und Navigieren zurück zur vorherigen Seite gelöscht. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

      Speichern Sie die Änderungen an **NoteEntryPage.xaml.cs** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

      > [!WARNING]
      > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

10. In der **Lösungspad**, wählen die **Anmerkungen zu dieser** -Projekt, mit der rechten Maustaste und wählen Sie **hinzufügen > neue Datei...** . In der **neue Datei** wählen Sie im Dialogfeld **Forms > Forms-ContentPage XAML**, nennen Sie die neue Datei **NotesPage**, und klicken Sie auf die **neu** Schaltfläche.

      Dadurch wird eine Seite namens hinzugefügt **NotesPage** in den Stammordner des Projekts. Auf dieser Seite werden die Stammseite der Anwendung.

11. In **NotesPage.xaml**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

    Dieser Code definiert deklarativ die Benutzeroberfläche auf der Seite besteht aus einem [ `ListView` ](xref:Xamarin.Forms.ListView) und [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem). Die `ListView` verwendet Daten binden, um Hinweise anzuzeigen, die von der Anwendung abgerufen werden, und wählen eine Notiz werden, navigieren Sie zu der `NoteEntryPage` , in dem die Anmerkung kann geändert werden kann. Alternativ kann eine neue Notiz erstellt werden, durch Drücken der `ToolbarItem`. Weitere Informationen zur Datenbindung finden Sie unter [Datenbindung](deepdive.md#data-binding) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

12. In **NotesPage.xaml.cs**, entfernen Sie den gesamten Vorlagencode, und Ersetzen Sie ihn durch den folgenden Code:

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

    Dieser Code definiert die Funktionalität für die `NotesPage`. Wenn die Seite wird angezeigt, die `OnAppearing` Methode ausgeführt wird, wird die füllt die [ `ListView` ](xref:Xamarin.Forms.ListView) mit Anmerkungen, die aus dem lokalen Anwendungsordner für Daten abgerufen wurden. Wenn die [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) gedrückt wird die `OnNoteAddedClicked` -Ereignishandler ausgeführt wird. Diese Methode navigiert zu der `NoteEntryPage`wird durch das Festlegen der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von der `NoteEntryPage` in ein neues `Note` Instanz. Wenn ein Element in der `ListView` ausgewählt ist die `OnListViewItemSelected` -Ereignishandler ausgeführt wird. Diese Methode navigiert zu der `NoteEntryPage`wird durch das Festlegen der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von der `NoteEntryPage` , die dem ausgewählten `Note` Instanz. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml.cs** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

    > [!WARNING]
    > Es wird versucht, die die Anwendung an diesem Punkt zu erstellen führen zu Fehlern, die in den nachfolgenden Schritten behoben werden.

13. In der **Lösungspad**, doppelklicken Sie auf **"App.Xaml.cs"** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
            ...
        }
    }
    ```

    Dieser Code Fügt eine Namespacedeklaration für die `System.IO` -Namespace, und fügt eine Deklaration für eine statische `FolderPath` Eigenschaft vom Typ `string`. Die `FolderPath` Eigenschaft wird verwendet, um den Pfad auf dem Gerät gespeichert, in denen sich Daten gespeichert werden. Darüber hinaus, dass der Code initialisiert die `FolderPath` -Eigenschaft in der `App` Konstruktor und initialisiert die [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) Eigenschaft eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) , hostet ein Instanz von `NotesPage`. Weitere Informationen zur Navigation finden Sie unter [Navigation](deepdive.md#navigation) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

14. In der **Lösungspad**in die **Anmerkungen zu dieser** Projekt, mit der rechten Maustaste **"MainPage.xaml"**, und wählen Sie **entfernen**. In der drücken Sie die angezeigten Dialogfeld die **löschen** Schaltfläche, um die Datei von der Festplatte zu entfernen.

    Dadurch wird eine Seite, die nicht mehr verwendet wird, entfernt.

15. Erstellen Sie und führen Sie das Projekt auf jeder Plattform. Weitere Informationen finden Sie unter [des Schnellstarts erstellen](single-page.md#building-the-quickstart).

    Auf der **NotesPage** drücken Sie die **+** Schaltfläche, um das Navigieren zu der **NoteEntryPage** und geben Sie einen Hinweis. Nach dem Speichern der Notiz. der Anwendungs navigiert zurück an die **NotesPage**.

    Geben Sie eine Anzahl von Anmerkungen zu dieser unterschiedlicher Länge, um das Verhalten der Anwendung zu beobachten.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In dieser schnellstartanleitung haben Sie gelernt, wie Sie:

- Fügen Sie zusätzliche Seiten zu einer Xamarin.Forms-Projektmappe hinzu.
- Führen Sie die Navigation zwischen Seiten.
- Verwenden Sie die Datenbindung zum Synchronisieren von Daten zwischen Elementen der Benutzeroberfläche und ihre Datenquelle ein.

Um die Anwendung zu ändern, damit sie ihre Daten in einer lokalen von SQLite.NET-Datenbank speichert, weiterhin mit dem nächsten Schnellstart fortfahren.

> [!div class="nextstepaction"]
> [Weiter](database.md)

## <a name="related-links"></a>Verwandte Links

- [Anmerkungen zu dieser Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)
- [Tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md)
