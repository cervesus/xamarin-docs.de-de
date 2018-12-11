---
title: Xamarin.Forms-Schnellstart
description: Dieser Artikel erklärt, wie Sie mit Xamarin.Forms eine einfache plattformübergreifende Notes-Anwendung erstellen, mit der Sie eine Notiz eingeben und auf dem Gerät speichern können.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/21/2018
ms.openlocfilehash: 880fa527d91098cf8c5f8c46cd9c5cce9ec9a489
ms.sourcegitcommit: 744c0a50420bb091fca8b92a84c20e61c741cf9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52743136"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms-Schnellstart

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)

Dieser Schnellstart erklärt, wie Sie mit Xamarin.Forms eine einfache plattformübergreifende Notes-Anwendung erstellen, mit der Sie eine Notiz eingeben und auf dem Gerät speichern können. Die fertige Anwendung wird unten gezeigt:

[![](quickstart-experimental-images/screenshots-sml.png "Notes-Anwendung")](quickstart-experimental-images/screenshots.png#lightbox "Notes-Anwendung")

::: zone pivot="windows"

## <a name="get-started-with-visual-studio"></a>Erste Schritte mit Visual Studio

1. Starten Sie Visual Studio auf dem **Startbildschirm**. Dadurch wird die Startseite geöffnet:

    ![](quickstart-experimental-images/vs/start-page.png "Visual Studio")

2. Klicken Sie in Visual Studio auf **Neues Projekt erstellen…**, um ein neues Projekt zu erstellen:

    ![](quickstart-experimental-images/vs/new-solution.png "Neues Projekt")

3. Klicken Sie im Dialogfeld **Neues Projekt** auf **Plattformübergreifend**, wählen Sie die Vorlage **Mobile App (Xamarin.Forms)** aus, geben Sie für „Name“ **Notes** ein, wählen Sie einen geeigneten Speicherort für das Projekt aus, und klicken Sie auf **OK**:

    ![](quickstart-experimental-images/vs/new-project.png "Plattformübergreifende Projektvorlagen")

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Schnellstart ist es erforderlich, dass die Projektmappe **Notes** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Schnellstart in die Projektmappe kopieren.

4. Klicken Sie im Dialogfeld **New Cross Platform App** (Neue plattformübergreifende App) auf **Leere App**, wählen Sie **.NET Standard** als Strategie für die Codefreigabe aus, und klicken Sie auf **OK**:

    ![](quickstart-experimental-images/vs/new-app.png "Neue plattformübergreifende App")

5. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notes** auf die Datei **MainPage.xaml**, um sie zu öffnen:

    ![](quickstart-experimental-images/vs/open-mainpage-xaml.png "„MainPage.xaml“ öffnen")

6. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="_editor"
                    Placeholder="Enter your note"
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

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Label`](xref:Xamarin.Forms.Label) zum Anzeigen von Text, einem [`Editor`](xref:Xamarin.Forms.Editor) für die Texteingabe und zwei [`Button`](xref:Xamarin.Forms.Button)-Instanzen besteht, die die Anwendung zum Speichern oder Löschen einer Datei anweisen. Die beiden `Button`-Instanzen sind horizontal in einem [`Grid`](xref:Xamarin.Forms.Grid) angeordnet, wobei die `Label`, `Editor` und `Grid` vertikal in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) angeordnet sind.

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

7. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **Notes**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen:

    ![](quickstart-experimental-images/vs/open-mainpage-codebehind.png "„MainPage.xaml.cs“ öffnen")

8. Entfernen Sie in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    Dieser Code definiert ein `_fileName`-Feld, das auf eine Datei namens `notes.txt` verweist, die Notes-Daten im lokalen Anwendungsdatenordner der Anwendung speichert. Wenn der Seitenkonstruktor ausgeführt wird, wird die Datei gelesen, falls sie vorhanden, und im [`Editor`](xref:Xamarin.Forms.Editor) angezeigt. Wenn die auf **Speichern** [`Button`](xref:Xamarin.Forms.Button) gedrückt haben, wird der `OnSaveButtonClicked`-Ereignishandler ausgeführt wird der Inhalt des `Editor` in die Datei gespeichert. Wenn Sie auf **Löschen**`Button` gedrückt haben, wird der `OnDeleteButtonClicked`-Ereignishandler ausgeführt, sodass die Datei (falls vorhanden) gelöscht und sämtlicher Text aus dem `Editor` entfernt wird.

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

### <a name="building-the-quickstart"></a>Erstellen des Schnellstarts

1. Klicken Sie in Visual Studio auf die Menüelemente **Erstellen > Projektmappe erstellen**, oder drücken Sie F6. Die Projektmappe wird erstellt, und eine Erfolgsmeldung wird in der Statusleiste von Visual Studio angezeigt:

      ![](quickstart-experimental-images/vs/build-succeeded.png "Erstellen erfolgreich")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Projektmappe erfolgreich erstellt wird.

2. Klicken Sie in der Symbolleiste von Visual Studio auf **Starten** (die dreieckige Wiedergabetaste), um die Anwendung in Ihrem ausgewählten Android-Emulator zu starten:

    ![](quickstart-experimental-images/vs/android-start.png "Android-Symbolleiste in Visual Studio")

    ![](quickstart-experimental-images/vs/notes-android.png "Notes im Android-Emulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

    > [!NOTE]
    > Die folgenden Schritte sollten nur durchgeführt werden, wenn Sie einen [gekoppelten Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) haben, der die Systemanforderungen für die Entwicklung von Xamarin.Forms erfüllt.

3. Klicken Sie in der Visual Studio-Symbolleiste mit der rechten Maustaste auf das Projekt **Notes.iOS**, und wählen Sie **Als Startprojekt festlegen** aus.

      ![](quickstart-experimental-images/vs/set-as-startup-project-ios.png "iOS als Startprojekt festlegen")

4. Drücken Sie in der Symbolleiste von Visual Studio die Schaltfläche **Start** (die dreieckige Schaltfläche, die einer Play-Taste ähnelt), um die Anwendung in dem von Ihnen gewählten [iOS-Remotesimulator](~/tools/ios-simulator/index.md) zu starten:

    ![](quickstart-experimental-images/vs/ios-start.png "iOS-Symbolleiste in Visual Studio")

    ![](quickstart-experimental-images/vs/notes-ios.png "Notes im iOS-Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

::: zone-end
::: zone pivot="macos"

## <a name="get-started-with-visual-studio-for-mac"></a>Erste Schritte mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und klicken Sie auf der Startseite auf **Neues Projekt...**, um ein neues Projekt zu erstellen:

    ![](quickstart-experimental-images/vsmac/new-project.png "Neue Projektmappe")

2. Klicken Sie im Dialogfeld **Vorlage für neues Projekt auswählen** auf **Multi-Plattform > App**, wählen Sie die Vorlage **Leere Forms-App** aus, und klicken Sie auf **Weiter**:

    ![](quickstart-experimental-images/vsmac/choose-template.png "Wählen einer Vorlage")

3. Benennen Sie im Dialog **Leere Formularanwendung konfigurieren** die neue Anwendung mit **Notes**, stellen Sie sicher, dass die Option **.NET-Standard verwenden** ausgewählt ist, und klicken Sie auf die Schaltfläche **Weiter**:    

    ![](quickstart-experimental-images/vsmac/configure-app.png "Forms-Anwendung konfigurieren")

4. Behalten Sie im Dialogfeld **Ihre neue leere Formularanwendung konfigurieren** die Namen für die Projektmappe und das Projekt als **Notes** bei, wählen Sie einen passenden Speicherort für das Projekt, und klicken Sie auf die Schaltfläche **Erstellen**, um das Projekt zu erstellen:

    ![](quickstart-experimental-images/vsmac/configure-project.png "Forms-Projekt konfigurieren")

    > [!IMPORTANT]
    > Die C#- und XAML-Codeausschnitte in diesem Schnellstart erfordern, dass die Projektmappe und das Projekt beide den Namen **Notes** tragen. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Schnellstart in das Projekt kopieren.

5. Doppelklicken Sie im **Lösungspad** im Projekt **Notes** auf die Datei **MainPage.xaml**, um sie zu öffnen:

    ![](quickstart-experimental-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="_editor"
                    Placeholder="Enter your note"
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

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Label`](xref:Xamarin.Forms.Label) zum Anzeigen von Text, einem [`Editor`](xref:Xamarin.Forms.Editor) für die Texteingabe und zwei [`Button`](xref:Xamarin.Forms.Button)-Instanzen besteht, die die Anwendung zum Speichern oder Löschen einer Datei anweisen. Die beiden `Button`-Instanzen sind horizontal in einem [`Grid`](xref:Xamarin.Forms.Grid) angeordnet, wobei die `Label`, `Editor` und `Grid` vertikal in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) angeordnet sind.

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

7. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **Notes**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen:

    ![](quickstart-experimental-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. Entfernen Sie in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    Dieser Code definiert ein `_fileName`-Feld, das auf eine Datei namens `notes.txt` verweist, die Notes-Daten im lokalen Anwendungsdatenordner der Anwendung speichert. Wenn der Seitenkonstruktor ausgeführt wird, wird die Datei gelesen, falls sie vorhanden, und im [`Editor`](xref:Xamarin.Forms.Editor) angezeigt. Wenn die auf **Speichern** [`Button`](xref:Xamarin.Forms.Button) gedrückt haben, wird der `OnSaveButtonClicked`-Ereignishandler ausgeführt wird der Inhalt des `Editor` in die Datei gespeichert. Wenn Sie auf **Löschen**`Button` gedrückt haben, wird der `OnDeleteButtonClicked`-Ereignishandler ausgeführt, sodass die Datei (falls vorhanden) gelöscht und sämtlicher Text aus dem `Editor` entfernt wird.

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

### <a name="building-the-quickstart"></a>Erstellen des Schnellstarts

1. Wählen Sie in Visual Studio für Mac die Menüelemente **Erstellen > Alle erstellen** aus, oder drücken Sie **&#8984;+B**. Das Projekt wird erstellt, und in der Symbolleiste von Visual Studio für Mac wird eine Erfolgsmeldung angezeigt.

      ![](quickstart-experimental-images/vsmac/build-successful.png "Buildvorgang erfolgreich")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis das Projekt erfolgreich erstellt wird.

2. Wählen Sie im **Lösungspad** das Projekt **Notes.iOS** aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Startprojekt festlegen** aus:

      ![](quickstart-experimental-images/vsmac/set-startup-project-ios.png "iOS als Startprojekt festlegen")

3. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator zu starten:

      ![](quickstart-experimental-images/vsmac/start.png "Symbolleiste in Visual Studio für Mac")

      ![](quickstart-experimental-images/vsmac/notes-ios.png "Notes im iOS-Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

4. Wählen Sie im **Lösungspad** das Projekt **Notes.Droid** aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Startprojekt festlegen** aus:

      ![](quickstart-experimental-images/vsmac/set-startup-project-android.png "Android als Startprojekt festlegen")

5. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten Android-Emulator zu starten:

      ![](quickstart-experimental-images/vsmac/notes-android.png "Notes im Android-Emulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

::: zone-end

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)
